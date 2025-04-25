using System;
using System.Reflection;
using MelonLoader;
using S1API.Items;

[assembly: MelonInfo(typeof(OGAssetsMod.OGAssets), "OG Assets", "1.0.0", "Sosa")]
[assembly: MelonGame("TVGS", "Schedule I")]

namespace OGAssetsMod
{
    public class OGAssets : MelonMod
    {
        public override void OnSceneWasLoaded(int buildIndex, string sceneName)
        {
            if (sceneName != "Main") return;
            try
            {
                MelonLogger.Msg("Attempting to rename 'viagor' item...");
                ItemDefinition viagorItem = ItemManager.GetItemDefinition("viagor");

                if (viagorItem != null)
                {
                    MelonLogger.Msg($"Found item: {viagorItem.ID}, current name: {viagorItem.Name}");
                    FieldInfo s1ItemDefField = typeof(ItemDefinition).GetField("S1ItemDefinition",
                        BindingFlags.NonPublic | BindingFlags.Instance);

                    if (s1ItemDefField != null)
                    {
                        var s1ItemDef = s1ItemDefField.GetValue(viagorItem);

                        if (s1ItemDef != null)
                        {
                            PropertyInfo nameProperty = s1ItemDef.GetType().GetProperty("Name");
                            if (nameProperty != null)
                            {
                                string oldName = (string)nameProperty.GetValue(s1ItemDef);
                                if (nameProperty.CanWrite)
                                {
                                    nameProperty.SetValue(s1ItemDef, "Viagra");
                                    MelonLogger.Msg($"Successfully renamed item from '{oldName}' to 'Viagra'.");
                                }
                                else
                                {
                                    MelonLogger.Warning("Name property is read-only. Trying to use a field instead...");
                                    FieldInfo nameField = s1ItemDef.GetType().GetField("_name",
                                        BindingFlags.NonPublic | BindingFlags.Instance);

                                    if (nameField != null)
                                    {
                                        nameField.SetValue(s1ItemDef, "Viagra");
                                        MelonLogger.Msg($"Successfully renamed item from '{oldName}' to 'Viagra' using field access.");
                                    }
                                    else
                                    {
                                        MelonLogger.Error("Could not find a way to modify the item name.");
                                    }
                                }
                            }
                        }
                        else
                        {
                            MelonLogger.Error("S1ItemDefinition is null.");
                        }
                    }
                    else
                    {
                        MelonLogger.Error("Could not access S1ItemDefinition field via reflection.");
                    }
                }
                else
                {
                    MelonLogger.Error("Failed to find item with ID 'viagor'.");
                }
            }
            catch (Exception ex)
            {
                MelonLogger.Error($"An error occurred: {ex}");
            }
        }
    }
}
